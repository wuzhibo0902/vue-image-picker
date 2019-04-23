# vue-image-picker
An image picker for image uploading.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run all tests
npm test
```

## Usage

### install by npm
```
npm install @wuzhibo/vue-image-picker --save
```

### script
```
// import
import ImagePicker from 'vue-image-picker'

// register as component
export default {
  //
  components: {
    ImagePicker
  },
  //
  methods: {
    // uploading by ajax
    ajaxUpload () {
      // get picked images
      const files = this.$refs['imagePicker].getFiles()
      //
      // TODO:...
      //
    }
  }
}
```

### template
```
<template>
  <div>
    <ImagePicker
      ref="imagePicker"
      :max="10">
  </div>
</template>
```

## Options

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| max | Number | 0 | The maximum of picked images. |
| multiple | Boolean | false | Enable support multiple pick |
| removeDuplication | Boolean | false | Enable remove duplicated images by file name |


## Methods

### getFiles()
get picked files, this return an array, such as:
```
[
  {
    title: 'filename', // filename for image title
    isCover: false, // is cover image
    file: file // H5 File Object
  },
  ...
]
```

### reset()
reset component status, typically used after upload successfully.


## license
MIT