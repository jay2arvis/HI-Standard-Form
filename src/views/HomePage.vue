<template>
  <ion-page>
    <multistep-form
        :fields="fields"
        :onFinish="onFinish"
        :onCancel="onCancel">
    </multistep-form>
  </ion-page>
</template>

<script lang="ts">
import { IonPage } from '@ionic/vue';
import { defineComponent } from 'vue';
import TextInput from "@/components/TextInput.vue"
import RadioSelect from "@/components/RadioSelect.vue"
import ItemList from "@/components/ItemList.vue"
import MultipleSelect from "@/components/MultiSelect.vue"
import BooleanSelect from "@/components/BooleanSelect.vue"
import MultistepForm from "@/forms/MultistepForm.vue"
import { FieldDataInterface, FieldInterface, Option } from '@/router/FieldInterfaces';

export default defineComponent({
  name: 'HomePage',
  components: {
    IonPage,
    MultistepForm
  },
  setup() {
    const fields: FieldInterface[] = [
      {
        id: 'first_name',
        helpText: 'First name',
        type: TextInput,
        isRequired: () => true
      },
      {
        id: 'last_name',
        helpText: 'Last name',
        type: TextInput,
        isRequired: () => true
      },
      {
        id: 'select_gender',
        helpText: 'Select Gender',
        type: RadioSelect,
        isRequired: () => true,
        options: () => {
          return [
            { label: 'Male', value: 'M'},
            { label: 'Female', value: 'F'}
          ]
        }
      },
      {
        id: 'multiple_select',
        helpText: 'Select hobbies',
        type: MultipleSelect,
        options: () => {
          return [
            { label: 'Sports', value: 'sports', isChecked: false},
            { label: 'Walking', value: 'Walking', isChecked: false},
            { label: 'Cycling', value: 'Cycling', isChecked: false},
            { label: 'Swimming', value: 'Swimming', isChecked: false}
          ]
        }
      },
      {
        id: 'truth_detector',
        helpText: 'Social history',
        type: BooleanSelect,
        options: () => {
          return [
            { label: "Smokes", value: '' },
            { label: "Drinks", value: '' },
            { label: "Married", value: '' }
          ]
        }
      },
      {
        id: 'summary',
        helpText: 'Summary',
        type: ItemList,
        options: (_, fieldData) => {
         const items: Option[] = []
         Object.values(fieldData).forEach((i: any) => {
          if (i.isAvailable && i.formValue != null) {
            if (Array.isArray(i.formValue)) {
              i.formValue.forEach((v: Option) => items.push({
                label: i.helpText,
                value: v.label
              }))
            } else {
              items.push({
                label: i.helpText,
                value: i.formValue.label
              })
            }
           }
         })
         return items
        },
        navButtons: {
          hide: ['Clear']
        }
      }
    ]
    function onFinish(data: Record<string, FieldDataInterface>) {
      console.log('onFinish', data)
    }
    function  onCancel() {
      console.log('Cancel button pressed!')
    }
    return {
      fields,
      onFinish,
      onCancel
    }
  }
});
</script>
