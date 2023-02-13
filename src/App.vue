<template>
  <div id="app" :style="backgroundStyle">
    <div class="flex">
      <background-picker @change="changeBackgroundColor" :label="colorLabel" :options="colorOptions" :pickerType="colorPickerType" :clearSelection="currentSelectionType != 'color'" size="sm"/>
      <background-picker @change="changeBackgroundImage" :label="imageLabel" :options="imageOptions" :pickerType="imagePickerType" :clearSelection="currentSelectionType != 'image'" size="md"/>
      <background-picker @change="changeBackgroundColor" :label="colorLabel" :options="colorOptions" :pickerType="colorPickerType" :clearSelection="currentSelectionType != 'color'" size="lg"/>
    </div>
    <div class="flex">
      <background-picker @change="changeBackgroundImage" :label="imageLabel" :options="imageOptions" :pickerType="imagePickerType" :clearSelection="currentSelectionType != 'image'" size="xl"/>
      <background-picker @change="changeBackgroundColor" :label="colorLabel" :options="colorOptions" :pickerType="colorPickerType" :clearSelection="currentSelectionType != 'color'" size="asd"/>
      <background-picker @change="changeBackgroundImage" :label="imageLabel" :options="imageOptions" :pickerType="imagePickerType" :clearSelection="currentSelectionType != 'image'" size="sm"/>
    </div>
  </div>
</template>

<script>
import BackgroundPicker from './components/BackgroundPicker.vue';
export default {
  name: "App",
  components: {BackgroundPicker},
  data() {
    return {
      backgroundStyle: "",
      colorLabel: "Color",
      colorOptions: ["#b0cfd2", "#e8bcbd", "#ddb0ca", "#edcc9a", "#e27978", "#b0cfd2", "#e8bcbd", "#ddb0ca"],
      colorPickerType: "color",
      imageLabel: "Imagen",
      imageOptions: [
        "https://images.pexels.com/photos/2832432/pexels-photo-2832432.png?auto=compress&cs=tinysrgb&dpr=1&w=500", 
        "https://img.freepik.com/free-vector/point-wave-noise-texture-abstract-dot-background-technological-cyberspace-background_1217-5375.jpg?w=2000", 
        "https://static-cse.canva.com/blob/560659/texture.jpg", 
        "https://cdn.shopify.com/s/files/1/0093/6227/6415/articles/238649ac73b8e600d3c4aca4ffeff237_640x.jpg?v=1609848102", 
        "https://mathvistu.files.wordpress.com/2017/02/soapfilm.jpg?w=1700",
        "https://free-3dtextureshd.com/wp-content/webpc-passthru.php?src=https://free-3dtextureshd.com/wp-content/uploads/2021/03/Gold-Textures-Seamless-square-wall-background-BPR-material-High-Resolution-Free-Download-HD-4k-preview-maps.jpg&nocache=1"
      ],
      imagePickerType: "image",
      currentSelectionType: ""
    }
  },
  methods: {
    changeBackgroundColor(item, itemType){
        this.backgroundStyle = "background-color:" + item + ";" ;
        this.currentSelectionType = "color"
    },
    changeBackgroundImage(item, itemType){
        this.backgroundStyle = "background-image: url(" + item + ");" ;
        this.currentSelectionType = "image"
    }
  }
}
</script>

<style>

</style>
