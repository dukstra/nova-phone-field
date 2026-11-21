<template>
    <DefaultField :field="field" :errors="errors" :show-help-text="showHelpText">
        <template #field>
            <input
                :id="field.name"
                type="text"
                ref="input"
                class="w-full form-control form-input form-input-bordered"
                :class="errorClasses"
                :placeholder="field.name"
                v-model="value"
            />
        </template>
    </DefaultField>
</template>

<script>
import PhoneMasks from '../data/phone-masks.json';
import Inputmask from 'inputmask';
import { FormField, HandlesValidationErrors } from 'laravel-nova';

export default {
    mixins: [FormField, HandlesValidationErrors],

    props: ['resourceName', 'resourceId', 'field'],

    data: () => ({
        builtInMasks: [],
        customMasks: []
    }),

    computed: {
        availableMasks() {
            let masks = this.builtInMasks.concat(this.customMasks);
            masks = masks.sort().map(mask => this.formatToInputmask(mask));

            return !masks.length ? '9{1,20}' : masks;
        }
    },

    mounted() {
        this.setBuiltInMasks();

        this.setCustomMasks();

        Inputmask(this.availableMasks).mask(this.$refs.input);
    },

    methods: {
        setInitialValue() {
            this.value = this.field.value || '';
        },

        setBuiltInMasks() {
            if (this.field.onlyCustomFormats) {
                this.builtInMasks = [];
                return;
            }

            let countries = this.field.onlyCountries || [];

            let masks = !countries.length ? PhoneMasks : this.loadMasksForCountries(countries);

            this.builtInMasks = masks.map(obj => obj.mask);
        },

        setCustomMasks() {
            this.customMasks = this.field.customFormats || [];
        },

        loadMasksForCountries(countries) {
            return PhoneMasks.filter(obj => countries.includes(obj.country_code));
        },

        formatToInputmask(value) {
            return value ? value.replace(/9/gi, '\\9').replace(/#/gi, '9') : '';
        },

        fill(formData) {
            formData.append(this.fieldAttribute, this.value || '');
        },

        handleChange(value) {
            this.value = value;
        }
    }
};
</script>
