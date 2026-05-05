# Chakra UI

## Table of contents

- [Chakra UI](#chakra-ui)
  - [Table of contents](#table-of-contents)
  - [What is ChakraUI?](#what-is-chakraui)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Theming and Customization](#theming-and-customization)
  - [Chakra UI Components](#chakra-ui-components)
  - [Component Examples](#component-examples)

## What is ChakraUI?

- An accessible UI component library
- A component library provides a bunch of component blocks that make developing a UI faster and more consistent.
- Available for both React and Vue

**Chakra UI** is a simple, modular, and accessible component library that provides the building blocks you need to build your React applications. It is a comprehensive library of accessible, reusable, and composable React components that streamlines the development of modern web applications and websites.

Here are some key features of Chakra UI:

- **Less Code, More Speed**: Spend less time writing UI code and more time building a great experience for your customers.
- **Accessible**: Chakra UI strictly follows WAI-ARIA standards for all components.
- **Themeable**: Customize any part of the components to match your design needs.
- **Composable**: Designed with composition in mind. Compose new components with ease.
- **Light and Dark UI**: Optimized for multiple color modes. Use light or dark, your choice.
- **Developer Experience**: Guaranteed to boost your productivity when building your app or website.
- **Active Community**: A team of active maintainers ready to help you whenever you need.

## Installation

- [Official Installation Guide](https://chakra-ui.com/getting-started/vite-guide)


  To install Chakra UI in your project, you can run the following commands in your terminal:

```bash
# Chakra UI v2 (React 18, Emotion-based)
npm i @chakra-ui/react @emotion/react @emotion/styled framer-motion

# Chakra UI v3 (2024 — new zero-config setup, no Emotion required)
npm i @chakra-ui/react
```

> **Version note:** **v3** (released 2024) is a full rewrite — no `@emotion` dependency, new `createSystem` theming API, and better performance. **v2** is still widely used. Check which version your project uses.

## Usage

After installing Chakra UI, you need to set up the `ChakraProvider` at the root of your application.

```jsx
import * as React from "react";
import { ChakraProvider } from "@chakra-ui/react";

function App() {
  return (
    <ChakraProvider>
      <TheRestOfYourApplication />
    </ChakraProvider>
  );
}
```

Please note that **v2** of Chakra UI is only compatible with React 18. **v3** supports React 18+.

## Theming and Customization

Chakra UI is fully themeable. Extend the default theme to match your brand:

```jsx
// v2 — extend the default theme
import { extendTheme, ChakraProvider } from '@chakra-ui/react';

const theme = extendTheme({
  colors: {
    brand: {
      50:  '#e3f9f5',
      500: '#319795',   // teal-500
      900: '#1a365d',
    },
  },
  fonts: {
    heading: `'Inter', sans-serif`,
    body:    `'Inter', sans-serif`,
  },
  config: {
    initialColorMode: 'light',   // 'light' | 'dark' | 'system'
    useSystemColorMode: false,
  },
});

function App() {
  return (
    <ChakraProvider theme={theme}>
      <YourApp />
    </ChakraProvider>
  );
}
```

```jsx
// Use brand colors anywhere via the `colorScheme` prop
<Button colorScheme="brand">Brand Button</Button>
```

### Dark Mode Toggle

```jsx
import { useColorMode, Button } from '@chakra-ui/react';

function DarkModeToggle() {
  const { colorMode, toggleColorMode } = useColorMode();
  return (
    <Button onClick={toggleColorMode}>
      Switch to {colorMode === 'light' ? 'Dark' : 'Light'} Mode
    </Button>
  );
}
```

## Chakra UI Components

- [Official Components Page](https://chakra-ui.com/docs/components)


  Chakra UI is a comprehensive library of accessible, reusable, and composable React components that streamlines the development of modern web applications and websites. It provides prebuilt components to help you build your React applications faster. Here are some of the component categories provided by Chakra UI:

1. **Layout**: Aspect Ratio, Box, Center, Container, Flex, Grid, SimpleGrid, Stack, Wrap
2. **Forms**: Button, Checkbox, Editable, Form Control, Icon Button, Input, Number Input, Pin Input, Radio, Range Slider, Select, Slider, Switch, Textarea
3. **Data Display**: Badge, Card, Code, Divider, Kbd, List, Stat, Table, Tag
4. **Feedback**: Alert, Circular Progress, Progress, Skeleton, Spinner, Toast
5. **Typography**: Text, Heading, Highlight
6. **Overlay**: Alert Dialog, Drawer, Menu, Modal, Popover, Tooltip
7. **Disclosure**: Accordion, Tabs, Visually Hidden
8. **Navigation**: Breadcrumb, Link, LinkOverlay, SkipNav, Stepper
9. **Media and Icons**: Avatar, Icon, Image
10. **Other**: Close Button, Portal, Show / Hide, Transitions

Each of these components is designed with accessibility in mind, adhering to WAI-ARIA standards. They are also themeable, allowing you to customize any part of the components to match your design needs.

## Component Examples

### Box, Flex, Stack

```jsx
import { Box, Flex, Stack, Text, Heading } from '@chakra-ui/react';

// Box = div with style props
<Box bg="gray.100" p={4} borderRadius="md">
  <Heading size="md">Card Title</Heading>
  <Text color="gray.600">Card body text.</Text>
</Box>

// Flex = flexbox container
<Flex justify="space-between" align="center">
  <Text>Left</Text>
  <Text>Right</Text>
</Flex>

// Stack = vertical spacing helper (HStack for horizontal)
<Stack spacing={3}>
  <Text>Item 1</Text>
  <Text>Item 2</Text>
  <Text>Item 3</Text>
</Stack>
```

### Form Controls

```jsx
import { FormControl, FormLabel, Input, FormErrorMessage, Button } from '@chakra-ui/react';

function EmailForm() {
  const [email, setEmail] = useState('');
  const isError = email === '';

  return (
    <FormControl isInvalid={isError}>
      <FormLabel>Email</FormLabel>
      <Input
        type="email"
        value={email}
        onChange={e => setEmail(e.target.value)}
        placeholder="Enter email"
      />
      {isError && <FormErrorMessage>Email is required.</FormErrorMessage>}
      <Button mt={4} colorScheme="teal" type="submit">Submit</Button>
    </FormControl>
  );
}
```

### Toast Notifications

```jsx
import { useToast, Button } from '@chakra-ui/react';

function NotifyButton() {
  const toast = useToast();
  return (
    <Button
      onClick={() =>
        toast({
          title: 'Saved!',
          description: 'Your changes have been saved.',
          status: 'success',   // 'success' | 'error' | 'warning' | 'info'
          duration: 3000,
          isClosable: true,
          position: 'top-right',
        })
      }
    >
      Save
    </Button>
  );
}
```

### Modal

```jsx
import { Modal, ModalOverlay, ModalContent, ModalHeader,
         ModalBody, ModalFooter, ModalCloseButton,
         useDisclosure, Button } from '@chakra-ui/react';

function DeleteModal() {
  const { isOpen, onOpen, onClose } = useDisclosure();

  return (
    <>
      <Button colorScheme="red" onClick={onOpen}>Delete</Button>
      <Modal isOpen={isOpen} onClose={onClose}>
        <ModalOverlay />
        <ModalContent>
          <ModalHeader>Confirm Delete</ModalHeader>
          <ModalCloseButton />
          <ModalBody>Are you sure? This action cannot be undone.</ModalBody>
          <ModalFooter>
            <Button variant="ghost" mr={3} onClick={onClose}>Cancel</Button>
            <Button colorScheme="red" onClick={() => { /* delete */ onClose(); }}>Delete</Button>
          </ModalFooter>
        </ModalContent>
      </Modal>
    </>
  );
}
```
