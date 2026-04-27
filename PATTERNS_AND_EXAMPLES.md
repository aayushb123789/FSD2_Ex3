# Responsive Design Patterns & Examples

This document provides detailed examples of responsive design patterns used throughout the project.

---

## 📱 Pattern 1: Mobile-First Grid System

### Basic 3-Column to 1-Column Layout

**Use Case:** Feature cards, product listings, team members

```jsx
<Grid container spacing={{ xs: 2, sm: 3, md: 4 }}>
  {items.map((item) => (
    <Grid item xs={12} sm={6} md={4} key={item.id}>
      <Card>
        <CardContent>
          {/* Card content */}
        </CardContent>
      </Card>
    </Grid>
  ))}
</Grid>
```

**Responsive Behavior:**
| Screen | Columns | Layout |
|--------|---------|--------|
| xs (0-599px) | 1 | Full width (12/12) |
| sm (600-959px) | 2 | Half width (6/12) |
| md (960px+) | 3 | One-third (4/12) |

**Key Points:**
- `xs={12}` - Takes full width on mobile
- `sm={6}` - Takes half width on tablets
- `md={4}` - Takes one-third on desktop
- Spacing increases with screen size

---

## 📱 Pattern 2: Hero Section with Responsive Typography

**Use Case:** Landing page banners, splash screens

```jsx
import { styled } from '@mui/material/styles';
import { Box, Container, Typography, Button } from '@mui/material';

const HeroBox = styled(Box)(({ theme }) => ({
  background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
  color: 'white',
  padding: theme.spacing(8, 0),
  [theme.breakpoints.down('md')]: {
    padding: theme.spacing(6, 0),
  },
  [theme.breakpoints.down('sm')]: {
    padding: theme.spacing(4, 0),
  },
}));

function HeroSection() {
  return (
    <HeroBox>
      <Container maxWidth="lg">
        <Typography
          variant="h2"
          sx={{
            fontSize: { xs: '2rem', sm: '2.5rem', md: '3rem' },
            fontWeight: 700,
            marginBottom: 2,
          }}
        >
          Welcome to Our Service
        </Typography>
        <Typography
          variant="h5"
          sx={{
            fontSize: { xs: '1rem', sm: '1.25rem', md: '1.5rem' },
            marginBottom: 4,
            opacity: 0.95,
          }}
        >
          Build beautiful, responsive interfaces with ease
        </Typography>
        <Box sx={{ display: 'flex', gap: 2, flexWrap: 'wrap' }}>
          <Button variant="contained">Get Started</Button>
          <Button variant="outlined">Learn More</Button>
        </Box>
      </Container>
    </HeroBox>
  );
}
```

**Key Techniques:**
- Typography scales at 3 breakpoints
- Padding adjusts based on screen size
- Container provides max-width
- Buttons wrap on small screens

---

## 📱 Pattern 3: Two-Column Layout (Stack on Mobile)

**Use Case:** Content + Sidebar, Image + Description

```jsx
<Container maxWidth="lg" sx={{ py: { xs: 4, md: 8 } }}>
  <Grid container spacing={4}>
    {/* Left Content */}
    <Grid
      item
      xs={12}
      md={6}
      sx={{
        display: 'flex',
        flexDirection: 'column',
        justifyContent: 'center',
      }}
    >
      <Typography variant="h3" sx={{ marginBottom: 2 }}>
        Main Content
      </Typography>
      <Typography color="textSecondary" paragraph>
        This content takes full width on mobile and half width on desktop.
      </Typography>
      <Button variant="contained" sx={{ width: 'fit-content' }}>
        Learn More
      </Button>
    </Grid>

    {/* Right Content */}
    <Grid item xs={12} md={6}>
      <Card sx={{ height: '100%', minHeight: 300 }}>
        <CardMedia
          component="img"
          image="/image.jpg"
          alt="Featured"
          sx={{ height: '100%', objectFit: 'cover' }}
        />
      </Card>
    </Grid>
  </Grid>
</Container>
```

**Responsive Behavior:**
- **Mobile (xs):** Content stacked vertically (full width)
- **Desktop (md):** Content side-by-side (50% each)
- Spacing grows with screen size

---

## 📱 Pattern 4: Responsive Navigation Bar

**Use Case:** Top navigation with mobile menu

```jsx
import { useState } from 'react';
import {
  AppBar,
  Toolbar,
  IconButton,
  Drawer,
  List,
  ListItem,
  useMediaQuery,
  useTheme,
  Box,
  Typography,
} from '@mui/material';
import MenuIcon from '@mui/icons-material/Menu';
import CloseIcon from '@mui/icons-material/Close';

function ResponsiveNavBar() {
  const [drawerOpen, setDrawerOpen] = useState(false);
  const theme = useTheme();
  const isMobile = useMediaQuery(theme.breakpoints.down('md'));

  const toggleDrawer = (open) => (event) => {
    if (
      event.type === 'keydown' &&
      (event.key === 'Tab' || event.key === 'Shift')
    ) {
      return;
    }
    setDrawerOpen(open);
  };

  const menuItems = ['Home', 'About', 'Services', 'Contact'];

  const drawerContent = (
    <List sx={{ width: 250 }}>
      {menuItems.map((item) => (
        <ListItem
          button
          key={item}
          onClick={() => setDrawerOpen(false)}
        >
          {item}
        </ListItem>
      ))}
    </List>
  );

  return (
    <AppBar position="static">
      <Toolbar>
        <Typography variant="h6" sx={{ flexGrow: 1 }}>
          Logo
        </Typography>

        {/* Desktop Navigation */}
        {!isMobile && (
          <Box sx={{ display: 'flex', gap: 3 }}>
            {menuItems.map((item) => (
              <Typography key={item} sx={{ cursor: 'pointer' }}>
                {item}
              </Typography>
            ))}
          </Box>
        )}

        {/* Mobile Hamburger */}
        {isMobile && (
          <IconButton
            color="inherit"
            onClick={toggleDrawer(true)}
          >
            {drawerOpen ? <CloseIcon /> : <MenuIcon />}
          </IconButton>
        )}

        {/* Mobile Drawer */}
        <Drawer
          anchor="right"
          open={drawerOpen}
          onClose={toggleDrawer(false)}
        >
          {drawerContent}
        </Drawer>
      </Toolbar>
    </AppBar>
  );
}
```

**Key Features:**
- Uses `useMediaQuery` to detect mobile
- Hamburger menu only on mobile (md down)
- Desktop navigation hidden on mobile
- Smooth drawer animation

---

## 📱 Pattern 5: Responsive Data Table

**Use Case:** User lists, admin panels

```jsx
import {
  Table,
  TableBody,
  TableCell,
  TableContainer,
  TableHead,
  TableRow,
  Paper,
} from '@mui/material';

function ResponsiveTable({ data }) {
  return (
    <TableContainer component={Paper}>
      <Table>
        <TableHead>
          <TableRow sx={{ backgroundColor: '#f5f5f5' }}>
            <TableCell>Name</TableCell>
            {/* Hide on mobile */}
            <TableCell sx={{ display: { xs: 'none', sm: 'table-cell' } }}>
              Email
            </TableCell>
            {/* Hide on mobile/tablet */}
            <TableCell sx={{ display: { xs: 'none', md: 'table-cell' } }}>
              Department
            </TableCell>
            <TableCell>Status</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {data.map((row) => (
            <TableRow key={row.id}>
              <TableCell>{row.name}</TableCell>
              <TableCell sx={{ display: { xs: 'none', sm: 'table-cell' } }}>
                {row.email}
              </TableCell>
              <TableCell sx={{ display: { xs: 'none', md: 'table-cell' } }}>
                {row.department}
              </TableCell>
              <TableCell>{row.status}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

**Responsive Behavior:**
- **Mobile:** Only Name, Status columns
- **Tablet:** Name, Email, Status
- **Desktop:** All columns visible

---

## 📱 Pattern 6: Responsive Sidebar Layout

**Use Case:** Dashboard, admin panel

```jsx
const DRAWER_WIDTH = 240;

function DashboardLayout() {
  const [drawerOpen, setDrawerOpen] = useState(false);
  const theme = useTheme();
  const isMobile = useMediaQuery(theme.breakpoints.down('md'));

  return (
    <Box sx={{ display: 'flex' }}>
      {/* Permanent Drawer (Desktop) */}
      {!isMobile && (
        <Drawer
          variant="permanent"
          sx={{
            width: DRAWER_WIDTH,
            flexShrink: 0,
            '& .MuiDrawer-paper': {
              width: DRAWER_WIDTH,
              boxSizing: 'border-box',
            },
          }}
        >
          {/* Sidebar Content */}
        </Drawer>
      )}

      {/* Temporary Drawer (Mobile) */}
      {isMobile && (
        <Drawer
          anchor="left"
          open={drawerOpen}
          onClose={() => setDrawerOpen(false)}
        >
          {/* Sidebar Content */}
        </Drawer>
      )}

      {/* Main Content */}
      <Box
        component="main"
        sx={{
          flexGrow: 1,
          width: isMobile ? '100%' : `calc(100% - ${DRAWER_WIDTH}px)`,
          padding: { xs: 2, sm: 3, md: 4 },
        }}
      >
        {/* Page Content */}
      </Box>
    </Box>
  );
}
```

**Key Features:**
- Desktop: Permanent sidebar
- Mobile: Hidden sidebar, toggleable drawer
- Main content adjusts width dynamically

---

## 📱 Pattern 7: Responsive Form Layout

**Use Case:** Contact forms, sign-up pages

```jsx
function ResponsiveForm() {
  return (
    <Container maxWidth="sm" sx={{ py: { xs: 4, md: 8 } }}>
      <Box
        component="form"
        sx={{
          display: 'grid',
          gridTemplateColumns: { xs: '1fr', sm: '1fr 1fr' },
          gap: { xs: 2, md: 3 },
        }}
      >
        {/* Full width field */}
        <TextField
          label="Full Name"
          fullWidth
          sx={{ gridColumn: { xs: '1', sm: '1 / -1' } }}
        />

        {/* Half width fields on desktop */}
        <TextField label="First Name" fullWidth />
        <TextField label="Last Name" fullWidth />

        {/* Full width field */}
        <TextField
          label="Email"
          fullWidth
          sx={{ gridColumn: { xs: '1', sm: '1 / -1' } }}
        />

        {/* Full width button */}
        <Button
          variant="contained"
          size="large"
          sx={{ gridColumn: { xs: '1', sm: '1 / -1' } }}
        >
          Submit
        </Button>
      </Box>
    </Container>
  );
}
```

---

## 📱 Pattern 8: Dark/Light Theme Toggle

**Use Case:** Theme switcher, user preferences

```jsx
import { useState } from 'react';
import { ThemeProvider, createTheme, CssBaseline } from '@mui/material';
import Brightness4Icon from '@mui/icons-material/Brightness4';
import Brightness7Icon from '@mui/icons-material/Brightness7';

function App() {
  const [darkMode, setDarkMode] = useState(false);

  const theme = createTheme({
    palette: {
      mode: darkMode ? 'dark' : 'light',
      primary: { main: '#667eea' },
      background: {
        default: darkMode ? '#121212' : '#ffffff',
      },
    },
  });

  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <Box sx={{ display: 'flex', justifyContent: 'flex-end', p: 2 }}>
        <IconButton
          onClick={() => setDarkMode(!darkMode)}
          color="inherit"
        >
          {darkMode ? <Brightness7Icon /> : <Brightness4Icon />}
        </IconButton>
      </Box>
      {/* Page content */}
    </ThemeProvider>
  );
}
```

---

## 📱 Pattern 9: Responsive Card Grid with Different Layouts

**Use Case:** Product showcase, portfolio grid

```jsx
<Grid container spacing={{ xs: 2, sm: 2, md: 3 }}>
  {products.map((product) => (
    <Grid
      item
      key={product.id}
      xs={12}     // Mobile: 1 column
      sm={6}      // Tablet: 2 columns
      md={4}      // Desktop: 3 columns
      lg={3}      // Large: 4 columns
    >
      <Card
        sx={{
          transition: 'all 0.3s',
          '&:hover': {
            transform: 'translateY(-8px)',
            boxShadow: 3,
          },
        }}
      >
        <CardMedia
          component="img"
          image={product.image}
          alt={product.name}
          sx={{
            height: { xs: 200, sm: 250, md: 300 },
            objectFit: 'cover',
          }}
        />
        <CardContent>
          <Typography variant="h6">{product.name}</Typography>
          <Typography variant="body2" color="textSecondary">
            {product.description}
          </Typography>
        </CardContent>
      </Card>
    </Grid>
  ))}
</Grid>
```

---

## 📱 Pattern 10: Responsive Modal/Dialog

**Use Case:** Confirmation dialogs, forms

```jsx
function ResponsiveDialog({ open, onClose }) {
  return (
    <Dialog
      open={open}
      onClose={onClose}
      maxWidth="sm"
      fullWidth
      fullScreen={{ xs: true, sm: false }}
      PaperProps={{
        sx: {
          m: { xs: 0, sm: 2 },
          maxWidth: { xs: '100%', sm: 500 },
        },
      }}
    >
      <DialogTitle>Dialog Title</DialogTitle>
      <DialogContent sx={{ p: { xs: 2, sm: 3 } }}>
        Content goes here
      </DialogContent>
      <DialogActions sx={{ gap: 1, p: { xs: 2, sm: 2 } }}>
        <Button onClick={onClose}>Cancel</Button>
        <Button variant="contained">Confirm</Button>
      </DialogActions>
    </Dialog>
  );
}
```

---

## 🎨 Advanced: Creating Responsive Styled Component

**Use Case:** Reusable components with responsive styles

```jsx
import { styled } from '@mui/material/styles';
import { Card, Box } from '@mui/material';

export const ResponsiveCard = styled(Card)(({ theme, variant = 'default' }) => ({
  borderRadius: 12,
  transition: 'all 0.3s ease-in-out',
  
  // Base styles
  backgroundColor: theme.palette.background.paper,
  padding: theme.spacing(3),
  
  // Responsive padding
  [theme.breakpoints.down('sm')]: {
    padding: theme.spacing(2),
  },
  
  // Responsive shadows
  boxShadow:
    variant === 'elevated'
      ? theme.shadows[8]
      : theme.shadows[2],
  
  // Hover effect
  '&:hover': {
    boxShadow: theme.shadows[12],
    transform: 'translateY(-4px)',
  },
}));

// Usage
<ResponsiveCard variant="elevated">
  Card Content
</ResponsiveCard>
```

---

## 📊 Responsive Comparison Table

| Pattern | Mobile | Tablet | Desktop | Use Case |
|---------|--------|--------|---------|----------|
| Grid Cards | 1 col | 2 col | 3-4 col | Products, features |
| Two Column | Stack | Stack | Side | Content + sidebar |
| Hero Section | Padded | Padded | Large | Landing pages |
| Navigation | Drawer | Drawer | Inline | Top nav bar |
| Data Table | Hide cols | Some cols | All cols | Admin panels |
| Sidebar | Hidden | Hidden | Visible | Dashboards |
| Form | Full | Half | Half | Data entry |
| Dialog | Fullscreen | Modal | Modal | Confirmations |

---

## 🚀 Performance Tips for Responsive Designs

1. **Use CSS-in-JS** (styled-components, @emotion)
   - Better performance than separate CSS files
   - Dynamic theming support

2. **Lazy Load Images**
   ```jsx
   <img loading="lazy" src="image.jpg" alt="..." />
   ```

3. **Optimize Media Queries**
   - Use `useMediaQuery` instead of CSS media queries
   - Prevent unnecessary re-renders

4. **CSS Grid vs Flexbox**
   - Grid: Complex multi-column layouts
   - Flexbox: Simple alignments

5. **Minimize DOM Nodes**
   - Avoid deeply nested components
   - Use CSS Grid instead of nested containers

---

This document serves as a reference guide for implementing responsive designs with Material UI. Each pattern includes working code and explanation of responsive behavior.
