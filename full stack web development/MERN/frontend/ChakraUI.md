# Table of contents:

- [Table of contents:](#table-of-contents)
- [Chakra UI](#chakra-ui)
  - [What is ChakraUI?](#what-is-chakraui)
  - [Installation](#installation)
  - [Usage](#usage)
  - [Chakra UI Components](#chakra-ui-components)

# Chakra UI

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
# For Vite
npm i @chakra-ui/react @emotion/react @emotion/styled framer-motion
```

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

Please note that version 2 of Chakra UI is only compatible with React 18.

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
