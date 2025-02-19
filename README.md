# Vue Hijri Calendar

A Vue.js library for displaying Hijri, Gregorian, and Umm al-Qura calendars.

## Features
- Display Hijri and Gregorian calendars
- Support for Umm al-Qura dates
- Easy to use and customize
- Lightweight and responsive design

## Installation
You can install the library via npm:

```bash
npm install vue-hijri-calendar
```

## Usage

### Global Registration
Register the component globally in your Vue application:

```javascript
import Vue from 'vue';
import VueHijriCalendar from 'vue-hijri-calendar';
import 'vue-hijri-calendar/dist/style.css'; // Import the CSS file

Vue.use(VueHijriCalendar);
```

### Local Registration
Use the component locally in your Vue component:

```javascript
import VueHijriCalendar from 'vue-hijri-calendar';
import 'vue-hijri-calendar/dist/style.css'; // Import the CSS file

export default {
  components: {
    VueHijriCalendar,
  },
};
```

### Example

```vue
<template>
  <div>
    <vue-hijri-calendar
      type="hijri" <!-- or 'gregorian' -->
      @date-selected="handleDateSelect"
    />
  </div>
</template>

<script>
export default {
  methods: {
    handleDateSelect(date) {
      console.log('Selected Date:', date);
    },
  },
};
</script>
```

## Props

| Prop Name | Type   | Default     | Description                       |
|-----------|--------|-------------|-----------------------------------|
| type      | String | 'gregorian' | Calendar type ('gregorian' or 'hijri') |

## Events

| Event Name    | Description                          |
|--------------|----------------------------------|
| date-selected | Emitted when a date is selected. |

## Building the Library
To build the library locally, follow these steps:

### Clone the repository:

```bash
git clone https://github.com/your-repo/vue-hijri-calendar.git
cd vue-hijri-calendar
```

### Install dependencies:

```bash
npm install
```

### Build the library:

```bash
npm run build
```

The build files will be generated in the `dist/` folder.

## Publishing to npm
To publish the library to npm, follow these steps:

1. Make sure you have an npm account. If not, sign up at [npmjs.com](https://www.npmjs.com/).
2. Log in to your npm account:

   ```bash
   npm login
   ```
3. Update the `version` field in `package.json` if needed.
4. Publish the package:

   ```bash
   npm publish
   ```

## Contributing
Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Commit your changes.
4. Submit a pull request.

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

