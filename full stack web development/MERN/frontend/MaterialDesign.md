# Material Design & MUI

## Table of contents

- [Material Design \& MUI](#material-design--mui)
  - [Table of contents](#table-of-contents)
  - [What is Material Design?](#what-is-material-design)
  - [Material Design 3 (M3) Core Principles](#material-design-3-m3-core-principles)
    - [Color System](#color-system)
    - [Typography Scale](#typography-scale)
    - [Elevation and Surface](#elevation-and-surface)
    - [Shape System](#shape-system)
  - [MUI (Material UI for React)](#mui-material-ui-for-react)
    - [Installation](#installation)
  - [Core Components](#core-components)
    - [Button](#button)
    - [TextField (Input)](#textfield-input)
    - [Card](#card)
    - [AppBar and Toolbar](#appbar-and-toolbar)
    - [Dialog (Modal)](#dialog-modal)
    - [Snackbar (Toast)](#snackbar-toast)
    - [Table](#table)
    - [Menu](#menu)
    - [Tabs](#tabs)
  - [Layout Components](#layout-components)
    - [Box](#box)
    - [Stack](#stack)
    - [Grid v2](#grid-v2)
    - [Container](#container)
  - [Theming](#theming)
    - [Creating a Theme](#creating-a-theme)
    - [Dark Mode](#dark-mode)
    - [CSS Variables Mode (MUI v6)](#css-variables-mode-mui-v6)
  - [The `sx` Prop](#the-sx-prop)
  - [Styled Components API](#styled-components-api)
  - [Icons](#icons)
  - [MUI v5 vs v6 — Key Changes](#mui-v5-vs-v6--key-changes)
  - [MUI vs Other Libraries](#mui-vs-other-libraries)
  - [Common Patterns](#common-patterns)
    - [Form with Validation](#form-with-validation)
    - [Responsive Drawer Navigation](#responsive-drawer-navigation)
  - [Common Pitfalls](#common-pitfalls)

---

## What is Material Design?

**Material Design** is Google's open-source design system — a set of guidelines, components, and tools for building high-quality digital experiences. It provides:

- A visual language based on real-world surfaces (paper, ink, light)
- Accessible, motion-rich interactive components
- A flexible theming system (color tokens, typography scale, shape)

**Versions:**
- **Material Design 1 (2014)** — original "card + FAB" aesthetic
- **Material Design 2 (2018)** — refined, expressive, used in Google apps
- **Material Design 3 / Material You (2021+)** — dynamic color, personalization, accessibility-first

**Official resource:** [m3.material.io](https://m3.material.io)

**MUI (Material UI)** is the most popular React implementation of Material Design (~90K GitHub stars).

---

## Material Design 3 (M3) Core Principles

### Color System

M3 uses a **tonal color palette** derived from a single seed color. It generates:

| Role | Usage |
|---|---|
| `primary` | Key actions, buttons, active states |
| `on-primary` | Text/icons on primary background |
| `primary-container` | Containers, filled tonal buttons |
| `secondary` | Supporting, less prominent components |
| `tertiary` | Contrasting accents |
| `error` | Error states |
| `surface` | Backgrounds, cards |
| `outline` | Borders, dividers |

Dynamic color (Material You) adapts the palette to the user's wallpaper on Android 12+.

### Typography Scale

M3 defines 15 type roles:

| Role | Size | Weight | Use Case |
|---|---|---|---|
| Display Large | 57px | Regular | Hero text |
| Display Medium | 45px | Regular | |
| Display Small | 36px | Regular | |
| Headline Large | 32px | Regular | Page titles |
| Headline Medium | 28px | Regular | |
| Headline Small | 24px | Regular | |
| Title Large | 22px | Regular | App bar title |
| Title Medium | 16px | Medium | |
| Title Small | 14px | Medium | |
| Label Large | 14px | Medium | Buttons |
| Label Medium | 12px | Medium | Chips, tabs |
| Label Small | 11px | Medium | |
| Body Large | 16px | Regular | Long-form text |
| Body Medium | 14px | Regular | |
| Body Small | 12px | Regular | Captions |

### Elevation and Surface

M3 uses **tonal elevation** (color tint) instead of heavy drop shadows:

- Higher surface → more primary color tint overlaid
- Level 0: no tint (background)
- Level 5: strongest tint (modal overlays)

### Shape System

M3 defines shape at component level: None → Extra Small → Small → Medium → Large → Extra Large → Full.

---

## MUI (Material UI for React)

### Installation

```bash
# Core + icons + date pickers
npm install @mui/material @emotion/react @emotion/styled
npm install @mui/icons-material
npm install @mui/x-date-pickers dayjs

# For MUI v6 CSS variables support
npm install @mui/material @mui/material-pigment-css
```

> **Peer dependency:** MUI uses [Emotion](https://emotion.sh/) for styling by default. You can swap to `styled-components` via the `styled-components` variant package.

Wrap your app with the provider:

```tsx
// main.tsx
import { ThemeProvider, CssBaseline } from '@mui/material';
import { theme } from './theme';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <ThemeProvider theme={theme}>
    <CssBaseline />   {/* normalize + apply background/text color from theme */}
    <App />
  </ThemeProvider>
);
```

---

## Core Components

### Button

```tsx
import Button from '@mui/material/Button';

// Variants
<Button variant="contained">Contained</Button>   // filled
<Button variant="outlined">Outlined</Button>     // bordered
<Button variant="text">Text</Button>             // no border/background

// Colors
<Button variant="contained" color="primary">Primary</Button>
<Button variant="contained" color="secondary">Secondary</Button>
<Button variant="contained" color="error">Delete</Button>
<Button variant="contained" color="success">Save</Button>

// Sizes
<Button size="small">Small</Button>
<Button size="medium">Medium</Button>
<Button size="large">Large</Button>

// With icon
import SendIcon from '@mui/icons-material/Send';
<Button variant="contained" endIcon={<SendIcon />}>Send</Button>

// Loading (MUI v6 / LoadingButton)
import { LoadingButton } from '@mui/lab';
<LoadingButton loading={isSubmitting} variant="contained">Submit</LoadingButton>
```

### TextField (Input)

```tsx
import TextField from '@mui/material/TextField';

// Basic
<TextField label="Email" type="email" fullWidth />

// Variants
<TextField variant="outlined" label="Outlined" />  // default
<TextField variant="filled"   label="Filled" />
<TextField variant="standard" label="Standard" />

// Validation / error state
<TextField
  label="Password"
  type="password"
  error={!!errors.password}
  helperText={errors.password?.message}
  {...register('password')}
/>

// Multiline
<TextField label="Description" multiline rows={4} fullWidth />

// Controlled
const [value, setValue] = useState('');
<TextField
  value={value}
  onChange={(e) => setValue(e.target.value)}
  label="Search"
  InputProps={{
    startAdornment: <InputAdornment position="start"><SearchIcon /></InputAdornment>
  }}
/>
```

### Card

```tsx
import { Card, CardContent, CardActions, CardMedia, CardHeader } from '@mui/material';

<Card sx={{ maxWidth: 345 }}>
  <CardMedia
    component="img"
    height="140"
    image="/product.jpg"
    alt="Product"
  />
  <CardHeader
    avatar={<Avatar>R</Avatar>}
    title="Card Title"
    subheader="Subtitle"
  />
  <CardContent>
    <Typography variant="body2" color="text.secondary">
      Card body content goes here.
    </Typography>
  </CardContent>
  <CardActions>
    <Button size="small">Learn More</Button>
    <Button size="small" color="primary">Share</Button>
  </CardActions>
</Card>
```

### AppBar and Toolbar

```tsx
import { AppBar, Toolbar, Typography, IconButton } from '@mui/material';
import MenuIcon from '@mui/icons-material/Menu';

<AppBar position="sticky" color="primary" elevation={1}>
  <Toolbar>
    <IconButton edge="start" color="inherit" onClick={toggleDrawer}>
      <MenuIcon />
    </IconButton>
    <Typography variant="h6" component="div" sx={{ flexGrow: 1 }}>
      My App
    </Typography>
    <Button color="inherit">Login</Button>
  </Toolbar>
</AppBar>
```

### Dialog (Modal)

```tsx
import { Dialog, DialogTitle, DialogContent, DialogContentText, DialogActions } from '@mui/material';

const [open, setOpen] = useState(false);

<Dialog open={open} onClose={() => setOpen(false)} maxWidth="sm" fullWidth>
  <DialogTitle>Confirm Delete</DialogTitle>
  <DialogContent>
    <DialogContentText>
      Are you sure you want to delete this item? This action cannot be undone.
    </DialogContentText>
  </DialogContent>
  <DialogActions>
    <Button onClick={() => setOpen(false)}>Cancel</Button>
    <Button onClick={handleDelete} color="error" variant="contained">
      Delete
    </Button>
  </DialogActions>
</Dialog>
```

### Snackbar (Toast)

```tsx
import { Snackbar, Alert } from '@mui/material';

const [open, setOpen] = useState(false);

<Snackbar
  open={open}
  autoHideDuration={4000}
  onClose={() => setOpen(false)}
  anchorOrigin={{ vertical: 'bottom', horizontal: 'center' }}
>
  <Alert severity="success" variant="filled" onClose={() => setOpen(false)}>
    Changes saved successfully!
  </Alert>
</Snackbar>
```

### Table

```tsx
import { Table, TableBody, TableCell, TableContainer, TableHead, TableRow, Paper } from '@mui/material';

<TableContainer component={Paper}>
  <Table sx={{ minWidth: 650 }}>
    <TableHead>
      <TableRow>
        <TableCell>Name</TableCell>
        <TableCell align="right">Age</TableCell>
        <TableCell align="right">Status</TableCell>
      </TableRow>
    </TableHead>
    <TableBody>
      {rows.map((row) => (
        <TableRow key={row.id} hover>
          <TableCell>{row.name}</TableCell>
          <TableCell align="right">{row.age}</TableCell>
          <TableCell align="right">
            <Chip label={row.status} color={row.status === 'active' ? 'success' : 'default'} size="small" />
          </TableCell>
        </TableRow>
      ))}
    </TableBody>
  </Table>
</TableContainer>
```

### Menu

```tsx
import { Menu, MenuItem, ListItemIcon, ListItemText } from '@mui/material';

const [anchorEl, setAnchorEl] = useState<null | HTMLElement>(null);

<IconButton onClick={(e) => setAnchorEl(e.currentTarget)}>
  <MoreVertIcon />
</IconButton>

<Menu anchorEl={anchorEl} open={Boolean(anchorEl)} onClose={() => setAnchorEl(null)}>
  <MenuItem onClick={handleEdit}>
    <ListItemIcon><EditIcon fontSize="small" /></ListItemIcon>
    <ListItemText>Edit</ListItemText>
  </MenuItem>
  <MenuItem onClick={handleDelete} sx={{ color: 'error.main' }}>
    <ListItemIcon><DeleteIcon fontSize="small" color="error" /></ListItemIcon>
    <ListItemText>Delete</ListItemText>
  </MenuItem>
</Menu>
```

### Tabs

```tsx
import { Tabs, Tab, Box } from '@mui/material';

const [tab, setTab] = useState(0);

<Box sx={{ borderBottom: 1, borderColor: 'divider' }}>
  <Tabs value={tab} onChange={(_, v) => setTab(v)}>
    <Tab label="Overview" />
    <Tab label="Details" />
    <Tab label="Reviews" />
  </Tabs>
</Box>

{tab === 0 && <OverviewPanel />}
{tab === 1 && <DetailsPanel />}
{tab === 2 && <ReviewsPanel />}
```

---

## Layout Components

### Box

`Box` is the fundamental layout component — a `div` with `sx` support.

```tsx
import Box from '@mui/material/Box';

<Box
  sx={{
    display: 'flex',
    alignItems: 'center',
    gap: 2,
    p: 3,
    bgcolor: 'background.paper',
    borderRadius: 2,
  }}
>
  Content
</Box>
```

### Stack

`Stack` is a flexbox container for 1D layouts with built-in spacing.

```tsx
import Stack from '@mui/material/Stack';

// Horizontal (row)
<Stack direction="row" spacing={2} alignItems="center">
  <Avatar /><Typography>John</Typography><Button>Follow</Button>
</Stack>

// Vertical (column) — default
<Stack spacing={3}>
  <TextField label="First Name" />
  <TextField label="Last Name" />
  <Button variant="contained">Submit</Button>
</Stack>

// Responsive direction
<Stack direction={{ xs: 'column', sm: 'row' }} spacing={2}>
```

### Grid v2

MUI v5.15+ ships Grid v2 (no more `item` / `container` props):

```tsx
import Grid from '@mui/material/Grid2';

<Grid container spacing={3}>
  <Grid size={12}>          {/* full width */}
    <Header />
  </Grid>
  <Grid size={{ xs: 12, md: 8 }}>   {/* responsive */}
    <MainContent />
  </Grid>
  <Grid size={{ xs: 12, md: 4 }}>
    <Sidebar />
  </Grid>
</Grid>
```

### Container

Centers content with max-width breakpoints:

```tsx
import Container from '@mui/material/Container';

<Container maxWidth="lg">      {/* lg = 1200px */}
  <App />
</Container>

// Full-width section
<Container disableGutters maxWidth={false}>
```

---

## Theming

### Creating a Theme

```tsx
// src/theme.ts
import { createTheme } from '@mui/material/styles';

export const theme = createTheme({
  palette: {
    mode: 'light',
    primary: {
      main:  '#6366f1',  // indigo
      light: '#818cf8',
      dark:  '#4f46e5',
    },
    secondary: {
      main: '#ec4899',   // pink
    },
    background: {
      default: '#f8fafc',
      paper:   '#ffffff',
    },
    text: {
      primary:   '#0f172a',
      secondary: '#64748b',
    },
  },
  typography: {
    fontFamily: '"Inter", "Roboto", sans-serif',
    h1: { fontSize: '2.25rem', fontWeight: 700 },
    h2: { fontSize: '1.875rem', fontWeight: 600 },
    button: { textTransform: 'none' },  // disable uppercase buttons
  },
  shape: {
    borderRadius: 8,
  },
  components: {
    // Override component defaults globally
    MuiButton: {
      defaultProps: { disableElevation: true },
      styleOverrides: {
        root: { borderRadius: '8px', fontWeight: 600 },
      },
    },
    MuiCard: {
      defaultProps: { elevation: 0 },
      styleOverrides: {
        root: { border: '1px solid', borderColor: '#e2e8f0' },
      },
    },
    MuiTextField: {
      defaultProps: { variant: 'outlined', size: 'small' },
    },
  },
});
```

### Dark Mode

```tsx
// Dynamic dark/light toggle
const [mode, setMode] = useState<'light' | 'dark'>('light');

const theme = useMemo(() =>
  createTheme({
    palette: { mode },
  }),
  [mode]
);

<ThemeProvider theme={theme}>
  <CssBaseline />
  <IconButton onClick={() => setMode(m => m === 'light' ? 'dark' : 'light')}>
    {mode === 'dark' ? <LightModeIcon /> : <DarkModeIcon />}
  </IconButton>
  <App />
</ThemeProvider>
```

### CSS Variables Mode (MUI v6)

MUI v6 supports CSS variables theming — enables SSR dark mode without flicker:

```tsx
import { createTheme, ThemeProvider } from '@mui/material/styles';

const theme = createTheme({ cssVariables: true });

<ThemeProvider theme={theme}>
  <App />
</ThemeProvider>
```

---

## The `sx` Prop

Every MUI component accepts an `sx` prop — an inline styling shorthand that maps to the theme.

```tsx
<Box sx={{
  // Theme-aware shortcuts
  p: 2,               // padding: theme.spacing(2) = 16px
  m: 1,               // margin: 8px
  bgcolor: 'primary.main',          // theme.palette.primary.main
  color: 'text.secondary',
  borderRadius: 2,    // theme.shape.borderRadius * 2 = 16px
  border: 1,          // 1px solid
  borderColor: 'divider',

  // Responsive
  fontSize: { xs: '0.875rem', md: '1rem' },
  display: { xs: 'none', sm: 'block' },

  // Pseudo-selectors
  '&:hover': { bgcolor: 'action.hover' },
  '& .MuiButton-root': { fontWeight: 700 },

  // Raw CSS fallback
  boxSizing: 'border-box',
}}>
```

**`sx` shorthand system:**

| `sx` key | CSS property | Example |
|---|---|---|
| `p`, `pt`, `pr`, `pb`, `pl`, `px`, `py` | padding | `p: 2` = 16px |
| `m`, `mt`, `mr`, `mb`, `ml`, `mx`, `my` | margin | `mt: 3` = 24px |
| `bgcolor` | background-color | `bgcolor: 'grey.100'` |
| `color` | color | `color: 'primary.main'` |
| `display` | display | `display: 'flex'` |
| `flexDirection`, `flexWrap`, `gap` | flex | `gap: 2` |
| `width`, `height`, `maxWidth`, `minHeight` | sizing | `width: '100%'` |
| `borderRadius` | border-radius | uses theme multiplier |
| `boxShadow` | box-shadow | `boxShadow: 3` |
| `typography` | all font props | `typography: 'h6'` |

---

## Styled Components API

For reusable styled MUI components with theme access:

```tsx
import { styled } from '@mui/material/styles';
import Card from '@mui/material/Card';

const StyledCard = styled(Card, {
  shouldForwardProp: (prop) => prop !== 'featured',
})<{ featured?: boolean }>(({ theme, featured }) => ({
  padding: theme.spacing(3),
  border: `1px solid ${featured ? theme.palette.primary.main : theme.palette.divider}`,
  backgroundColor: featured ? theme.palette.primary.light : theme.palette.background.paper,
  transition: theme.transitions.create(['border-color', 'box-shadow']),
  '&:hover': {
    boxShadow: theme.shadows[4],
  },
}));

// Usage
<StyledCard featured>Featured content</StyledCard>
```

---

## Icons

```bash
npm install @mui/icons-material
```

```tsx
import HomeIcon        from '@mui/icons-material/Home';
import DeleteIcon      from '@mui/icons-material/Delete';
import FavoriteIcon    from '@mui/icons-material/Favorite';
import CheckCircleIcon from '@mui/icons-material/CheckCircle';

// Size
<HomeIcon fontSize="small" />   // 20px
<HomeIcon fontSize="medium" />  // 24px (default)
<HomeIcon fontSize="large" />   // 35px

// Color
<DeleteIcon color="error" />
<CheckCircleIcon color="success" />
<FavoriteIcon color="primary" />

// Custom size/color via sx
<HomeIcon sx={{ fontSize: 40, color: 'primary.main' }} />
```

Browse all icons at [mui.com/material-ui/material-icons](https://mui.com/material-ui/material-icons/).

---

## MUI v5 vs v6 — Key Changes

| Feature | v5 | v6 |
|---|---|---|
| Grid | `<Grid item xs={6}>` | `<Grid size={6}>` (Grid v2) |
| CSS Variables | Not supported | `cssVariables: true` in theme |
| Dark mode SSR | Flicker possible | Flicker-free with CSS vars |
| Emotion version | Emotion 11 | Emotion 11 (same) |
| Bundle size | ~100KB gzipped | Slightly smaller (tree-shaking improvements) |
| `sx` performance | Re-creates on every render | Improved caching |
| `LoadingButton` | `@mui/lab` | Still in `@mui/lab` |
| Pigment CSS | Not available | Optional zero-runtime CSS-in-JS |

---

## MUI vs Other Libraries

| Library | Philosophy | Bundle | Learning Curve | Best For |
|---|---|---|---|---|
| **MUI** | Material Design spec, comprehensive | ~100KB | Medium | Dashboards, admin UIs, enterprise apps |
| **Chakra UI** | Accessible, composable, minimal opinions | ~60KB | Low | React apps needing flexibility |
| **Tailwind + shadcn/ui** | Utility-first + headless | Pay-per-class | Medium | Full design control |
| **Ant Design** | Enterprise design system | ~150KB | Medium-High | B2B, data-heavy interfaces |
| **Radix UI** | Headless primitives, zero styles | Minimal | Low-Medium | Custom design systems |
| **Bootstrap** | CSS-first, broad browser support | ~60KB | Low | Quick prototypes, non-React |

---

## Common Patterns

### Form with Validation

```tsx
import { useForm, Controller } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const schema = z.object({
  email:    z.string().email(),
  password: z.string().min(8),
});

type FormData = z.infer<typeof schema>;

function LoginForm() {
  const { control, handleSubmit, formState: { errors, isSubmitting } } = useForm<FormData>({
    resolver: zodResolver(schema),
  });

  return (
    <Box component="form" onSubmit={handleSubmit(onSubmit)}>
      <Stack spacing={2}>
        <Controller name="email" control={control} render={({ field }) => (
          <TextField
            {...field}
            label="Email"
            type="email"
            fullWidth
            error={!!errors.email}
            helperText={errors.email?.message}
          />
        )} />
        <Controller name="password" control={control} render={({ field }) => (
          <TextField
            {...field}
            label="Password"
            type="password"
            fullWidth
            error={!!errors.password}
            helperText={errors.password?.message}
          />
        )} />
        <LoadingButton loading={isSubmitting} type="submit" variant="contained" fullWidth>
          Sign In
        </LoadingButton>
      </Stack>
    </Box>
  );
}
```

### Responsive Drawer Navigation

```tsx
const DRAWER_WIDTH = 240;

function Layout({ children }) {
  const [mobileOpen, setMobileOpen] = useState(false);
  const isMobile = useMediaQuery(theme.breakpoints.down('md'));

  const drawer = (
    <Box>
      <Toolbar />
      <List>
        {navItems.map((item) => (
          <ListItem key={item.text} disablePadding>
            <ListItemButton selected={pathname === item.href} component={Link} href={item.href}>
              <ListItemIcon>{item.icon}</ListItemIcon>
              <ListItemText primary={item.text} />
            </ListItemButton>
          </ListItem>
        ))}
      </List>
    </Box>
  );

  return (
    <Box sx={{ display: 'flex' }}>
      <AppBar position="fixed" sx={{ zIndex: (t) => t.zIndex.drawer + 1 }}>
        <Toolbar>
          {isMobile && (
            <IconButton color="inherit" edge="start" onClick={() => setMobileOpen(true)}>
              <MenuIcon />
            </IconButton>
          )}
          <Typography variant="h6" noWrap>My App</Typography>
        </Toolbar>
      </AppBar>

      {/* Mobile drawer */}
      <Drawer variant="temporary" open={mobileOpen} onClose={() => setMobileOpen(false)}
        ModalProps={{ keepMounted: true }}
        sx={{ display: { xs: 'block', md: 'none' }, '& .MuiDrawer-paper': { width: DRAWER_WIDTH } }}>
        {drawer}
      </Drawer>

      {/* Desktop drawer */}
      <Drawer variant="permanent"
        sx={{ display: { xs: 'none', md: 'block' }, '& .MuiDrawer-paper': { width: DRAWER_WIDTH } }}
        open>
        {drawer}
      </Drawer>

      <Box component="main" sx={{ flexGrow: 1, p: 3, mt: 8, ml: { md: `${DRAWER_WIDTH}px` } }}>
        {children}
      </Box>
    </Box>
  );
}
```

---

## Common Pitfalls

| Problem | Cause | Fix |
|---|---|---|
| `sx` spacing not matching design | `p: 2` = 16px, not 2px | Remember: 1 unit = 8px by default |
| Button text is UPPERCASE | Default MUI style | Add `typography: { button: { textTransform: 'none' } }` in theme |
| Dark mode flash on SSR | Theme not applied server-side | Use `cssVariables: true` (MUI v6) or `InitColorSchemeScript` |
| Theme not applied to Portal components (Menu, Dialog) | Portal renders outside ThemeProvider subtree | Portals automatically inherit theme in MUI — if not, check nesting |
| Large bundle | Importing from index | Use named imports: `import Button from '@mui/material/Button'` not `import { Button } from '@mui/material'` — both tree-shake in modern bundlers, but named is more explicit |
| `ref` forwarding error | Wrapping MUI component without forwardRef | Use `Box component={CustomComponent}` or ensure custom component forwards ref |
