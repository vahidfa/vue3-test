<template>
  <Dialog
    v-model:visible="show"
    :style="{ width: '600px' }"
    header="فرم"
    :modal="true"
    class="p-fluid"
  >
    <div v-for="item of temp" class="p-field" :key="item">
      <div v-if="item?.type === 'text'">
        <label :for="item.label">{{ item.label }}</label>
        <InputText
          :id="item.label"
          v-model="item.selected"
          :placeholder="item.placeholder"
          :required="item.require"
        />
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >این فیلد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'password'">
        <label :for="item.label">{{ item.label }}</label>
        <Password
          :id="item.label"
          v-model="item.selected"
          :placeholder="item.placeholder"
          :required="item.require"
        />
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >این فیلد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'number'">
        <label :for="item.label">{{ item.label }}</label>
        <InputNumber
          :id="item.label"
          v-model="item.selected"
          :required="item.require"
        />
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >این فیلد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'textarea'">
        <label :for="item.label">{{ item.label }}</label>
        <Textarea
          :id="item.label"
          v-model="item.selected"
          :required="item.require"
          rows="3"
          cols="20"
        />
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >این فیلد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'select'">
        <label :for="item.label" class="p-mb-3">{{ item.label }}</label>
        <Dropdown
          :id="item.label"
          v-model="item.selected"
          :options="item.items"
          optionLabel="label"
          :placeholder="item.placeholder"
        />
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >این فیلد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'multiselect'">
        <label :for="item.label" class="p-mb-3">{{ item.label }}</label>
        <MultiSelect
          :id="item.label"
          v-model="item.selected"
          :options="item.items"
          optionLabel="label"
          :placeholder="item.placeholder"
        />
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >این فیلد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'radiobutton'">
        <label>{{ item.label }}</label>
        <div v-for="radio in item.items" :key="radio">
          <RadioButton
            :id="radio.label"
            :name="radio.label"
            :value="radio"
            v-model="item.selected"
          />
          <label :for="radio.label">{{ radio.label }}</label>
        </div>
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >انتخاب یک مورد الزامی است</span
        >
      </div>
      <div v-if="item?.type === 'checkbox'">
        <label>{{ item.label }}</label>
        <div v-for="check in item.items" :key="check">
          <Checkbox
            :id="check.label"
            :name="check.label"
            :value="check"
            v-model="item.selected"
          />
          <label :for="check.label">{{ check.label }}</label>
        </div>
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >انتخاب یک یا چند مورد الزامی است.</span
        >
      </div>
      <div v-if="item?.type === 'file'">
        <label>{{ item.label }}</label>
        <FileUpload
          :name="item.name"
          :url="item.url"
          @upload="onUpload"
          :multiple="(item as IFile).multiple"
          :accept="item.accept"
          chooseLabel="انتخاب فایل"
          uploadLabel="آپلود"
          cancelLabel="انصراف"
          :maxFileSize="item.maxFileSize || 1000"
        >
          <template #empty>
            <p>با کشیدن و رها کردن فایل را آپلود کنید.</p>
          </template>
        </FileUpload>
        <span class="p-error" v-if="submitted && !item.selected && item.require"
          >انتخاب فایل الزامی است</span
        >
      </div>
    </div>
    <template #footer>
      <Button
        icon="pi pi-times"
        class="p-button-rounded p-button-danger p-button-outlined"
        @click="show = false"
      />
      <Button
        icon="pi pi-check"
        class="p-button-rounded p-button-outlined"
        @click="formSubmit"
      />
    </template>
  </Dialog>
  <Button @click="show = true">show</Button>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";
import Dialog from "primevue/dialog";
import InputText from "primevue/inputtext";
import Textarea from "primevue/textarea";
import InputNumber from "primevue/inputnumber";
import RadioButton from "primevue/radiobutton";
import Dropdown from "primevue/dropdown";
import MultiSelect from "primevue/multiselect";
import Checkbox from "primevue/checkbox";
import FileUpload from "primevue/fileupload";
import Button from "primevue/button";
import Password from "primevue/password";

interface IInputText {
  label: string;
  placeholder: string;
  type: string;
  require?: boolean;
  selected: string;
}
interface IInputPassword {
  label: string;
  placeholder: string;
  type: string;
  require?: boolean;
  selected: string;
}

interface IInputNumber {
  label: string;
  placeholder: string;
  type: string;
  require?: boolean;
  selected: number;
}

interface ITextArea {
  label: string;
  placeholder: string;
  type: string;
  require?: boolean;
  selected: string;
}
interface ISelect {
  label: string;
  placeholder: string;
  type: string;
  require?: boolean;
  selected: {
    value: string | boolean | number;
    label: string;
  };
  items: [
    {
      value: string | boolean | number;
      label: string;
    }
  ];
}

interface IMultiSelect {
  label: string;
  placeholder: string;
  type: string;
  require?: boolean;
  selected: [
    {
      value: string | boolean | number;
      label: string;
    }
  ];
  items: [
    {
      value: string | boolean | number;
      label: string;
    }
  ];
}

interface IRadioButton {
  label: string;
  type: string;
  require?: boolean;
  selected: {
    value: string | boolean | number;
    label: string;
  };
  items: [
    {
      value: string | boolean | number;
      label: string;
    }
  ];
}
interface ICheckBox {
  label: string;
  type: string;
  require?: boolean;
  selected: {
    value: string | boolean | number;
    label: string;
  };
  items: [
    {
      value: string | boolean | number;
      label: string;
    }
  ];
}

interface IFile {
  name: string;
  require?: boolean;
  url: string;
  type: string;
  label: string;
  maxFileSize: null;
  multiple: boolean;
  accept: string;
}

interface ITemp {
  text?: IInputText;
  password: IInputPassword;
  number?: IInputNumber;
  textarea?: ITextArea;
  select?: ISelect;
  multiselect?: IMultiSelect;
  radiobutton?: IRadioButton;
  checkbox?: ICheckBox;
  file?: IFile;
}

type Data =
  | IInputText["selected"]
  | IInputNumber["selected"]
  | ITextArea["selected"]
  | ISelect["selected"]
  | IMultiSelect["selected"]
  | IRadioButton["selected"]
  | ICheckBox["selected"];

export default defineComponent({
  components: {
    Dialog,
    InputText,
    InputNumber,
    Textarea,
    RadioButton,
    Dropdown,
    MultiSelect,
    Checkbox,
    FileUpload,
    Button,
    Password,
  },
  props: {
    model: {
      type: Object as PropType<ITemp>,
      default: () => ({}),
    },
    data: Object,
  },
  data() {
    return {
      values: {},
      show: false,
      temp: {} as ITemp,
      submitted: false,
    };
  },
  mounted() {
    this.temp = this.model;
  },
  methods: {
    formSubmit() {
      this.submitted = true;
      const data: Record<string, Data> = {};
      for (const key in this.temp) {
        data[key] = (this.temp[key as keyof ITemp] as any)?.selected;
      }
      this.$emit("data", data);
    },
    onUpload() {
      return false;
    },
  },
});
</script>

<style scoped>
* {
  direction: rtl;
}
.p-button {
  width: 36px !important;
}
</style>
