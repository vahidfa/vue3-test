<template>
  <div class="home">
    <FormGenerator :model="model" @data="getData" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import FormGenerator from "@/components/FormGenerator.vue";
export default defineComponent({
  name: "Home",
  components: {
    FormGenerator,
  },
  data() {
    return {
      data: {},
      model: {
        name: {
          label: "نام",
          placeholder: "نام",
          type: "text",
          require: true,
          selected: "",
        },
        age: {
          label: "سن",
          placeholder: "سن",
          type: "number",
          require: false,
          selected: "",
        },
        description: {
          label: "توضیحات",
          placeholder: "توضیحات",
          type: "textarea",
          require: true,
          selected: "",
        },
        country: {
          label: "کشور",
          placeholder: "کشور",
          type: "select",
          require: true,
          selected: "",
          items: [
            {
              value: "ایران",
              label: "ایران",
            },
            {
              value: "برزیل",
              label: "برزیل",
            },
          ],
        },
        city: {
          label: "شهر",
          placeholder: "انتخاب شهر",
          type: "multiselect",
          selected: "",
          require: true,
          items: [
            {
              value: "shiraz",
              label: "shiraz",
            },
            {
              value: "tehran",
              label: "tehran",
            },
          ],
        },
        category: {
          label: "دسته بندی",
          type: "radiobutton",
          require: true,
          selected: "",
          items: [
            {
              value: "general",
              label: "general",
            },
            {
              value: "expert",
              label: "expert",
            },
          ],
        },
        tags: {
          label: "برچسب ها",
          type: "checkbox",
          selected: "",
          require: true,
          items: [
            {
              value: "movie",
              label: "فیلم",
            },
            {
              value: "serial",
              label: "سریال",
            },
            {
              value: "animation",
              label: "انیمیشن",
            },
          ],
        },
        // image?: {
        //   name: string;
        //   url: string;
        //   type: string;
        //   label: string;
        //   maxFileSize: null;
        //   multiple: boolean;
        //   accept: string;
        // };
      },
    };
  },
  methods: {
    getData(data: any) {
      this.data = data;
    },
  },
});
</script>
