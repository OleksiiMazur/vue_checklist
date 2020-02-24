<template>
    <div class="main-wrapp">
        <div class="title-group">
            <h1>
                {{ title }}
            </h1>
            <p class="quantity">
                Now You have
                <span v-if="checklist.length > 0">{{ checklist.length }}</span>
                <span v-else>no</span> tasks.
            </p>
        </div>

        <form @submit.prevent="addItem">
            <input
                type="text"
                placeholder="Start typing here..."
                name="listItem"
                v-model="listItem"
                :class="{errored: ($v.listItem.$dirty && !$v.listItem.required) || ($v.listItem.$dirty && !$v.listItem.minLength)}"
            >
            <div class="params">
                <p v-if="($v.listItem.$dirty && !$v.listItem.required) || ($v.listItem.$dirty && !$v.listItem.minLength)"
                    class="form__help-block">
                        Type at least {{$v.listItem.$params.minLength.min}} characters
                </p>
                <div class="colors-wrap">
                    <div class="colors">
                        <span
                            v-for="color in colors"
                            :key="color.colorName">
                                <input
                                    :id="color.colorName"
                                    type="radio"
                                    v-bind:value="color.colorName"
                                    v-model="colorType"
                                    :name="colorType.index"
                                    required="required">
                                <label
                                    v-bind:class="color.colorName"
                                    :for="color.colorName"
                                    :key="color.colorName">
                                </label>
                        </span>
                    </div>
                    <button
                        type="submit"
                        v-bind:click.prevent="addItem">
                        Create
                    </button>
                    <p
                        v-if="($v.colorType.$dirty && !$v.colorType.required)"
                        class="form__help-block">
                        Choose a color, pleace 🌈
                    </p>
                </div>
            </div>
        </form>
        
        <p v-if="checklist.length < 1"
            class="create-new">
            Create a new item
        </p>
        <ul v-else class="checklist">
            <li
                :class="'checklist__item ' + data.itemColor"
                v-for="(data, index) in checklist"
                :key="index">
                    <p>
                        {{ data.itemName }}
                        <span v-on:click="removeItem">
                            x
                        </span>
                    </p>
            </li>
            <li v-if="listItem.length > 0"
                :class="'checklist__item checklist__item--new-item ' + colorType">
                    {{ listItem }}
            </li>
        </ul>
</div>
</template>

<script>
    import {required, minLength} from 'vuelidate/lib/validators'
    
    export default {
        name: 'Checklist',
        props: {},
        data() {
            return {
                title: 'Checklist',
                colors: [
                    {'colorName': 'green'},
                    {'colorName': 'orange'},
                    {'colorName': 'red'},
                    {'colorName': 'black'},
                    {'colorName': 'white'},
                ],
                checklist: [
                    {
                        'itemName': 'first',
                        'itemColor': 'white'
                    },
                    {
                        'itemName': 'second',
                        'itemColor': 'white'
                    }
                ],
                listItem: '',
                colorType: '',
            }
        },
        validations: {
            listItem: {
                required,
                minLength: minLength(4),
            },
            colorType: {
                required,
            },
        },
        watch: {},
        computed: {},
        methods: {
            addItem() {
                if (this.$v.$invalid) {
                    this.$v.$touch();
                    return
                }
                const checklistFormItem = {
                    listItem: this.listItem,
                    colorType: this.colorType,
                };

                this.checklist.push({itemName: this.listItem, itemColor: this.colorType});
                this.listItem = '';
                this.colorType = '';
                this.$v.$reset();
                
                console.log(checklistFormItem);
            },
            removeItem(id) {
                console.log(this.listItem.index);
                this.checklist.splice(this.checklist.id, 1);
            }
        },
    }
</script>

<style lang="scss">
    @import '../assets/css/checklist-default';
</style>
