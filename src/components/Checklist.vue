<template>
    <div class="main-wrapp">
        <h1>
            {{ title }}
        </h1>

        <form @submit.prevent="addItem">
            <input
                type="text"
                placeholder="Start typing here..."
                v-validate="'min:4'"
                name="listItem"
                v-model="listItem">
            
            <div class="params">
                <p class="form__help-block"
                   v-if="errors.has('listItem')">
                        Type at least 4 characters
                </p>
                <div v-else-if="listItem.length > 3"
                     class="colors-wrap">
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
                                    v-validate="'required|included:' + colorType"
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
                </div>
                <p v-else-if="errors.has('colorType')">
                    Choose a color, pleace 🌈🖌🖍✏
                </p>
                <p v-else
                    class="form__help-block quantity">
                        Now You have <span>{{ checklist.length }}</span> tasks.
                </p>
            </div>
        </form>
        
        <p v-if="checklist.length < 1">
            Create a new item
        </p>
        <ul v-else class="checklist">
            <li
                :class="'checklist__item ' + data.itemColor"
                v-for="(data, index) in checklist"
                :key="index">
                    {{ data.itemName }}
            </li>
            <li v-if="listItem.length > 0"
                :class="'checklist__item checklist__item--new-item ' + colorType">
                    {{ listItem }}
            </li>
        </ul>
</div>
</template>

<script>
    import { required, minLength, maxLength, between, email } from 'vee-validate'
    
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
                listItem: '',
                colorType: '',
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
            }
        },
        methods: {
            addItem() {
                this.$validator.validateAll()
                    .then((result) => {
                        if (result && this.listItem.length > 3) {
                            this.checklist.push({itemName: this.listItem, itemColor: this.colorType});
                            this.listItem = '';
                        } else if (this.listItem.length < 4) {
                            console.log('At least 4 characters');
                        } else {
                            console.log('Chose a color');
                        }
                    });
            }
        },
    }
</script>

<style lang="scss">
    @import '../assets/css/checklist-default';
</style>
